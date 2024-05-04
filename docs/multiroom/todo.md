# Reste à faire

## Autorisation Bluetooth

Pour le moment, à chaque connexion, une autorisation est nécessaire

``` shell
pi@multiroom1:~ $ bluetoothctl
Agent registered
[CHG] Controller B8:27:EB:5C:D0:BF Pairable: yes
[CHG] Device 74:B5:87:92:0D:35 Connected: yes
Authorize service
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 00000000-deca-fade-deca-deafdecacafe
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 00001000-0000-1000-8000-00805f9b34fb
...
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 0000180f-0000-1000-8000-00805f9b34fb
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 02030302-1d19-415f-86f2-22a2106a0a77
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 1ff31936-572e-4b36-a2bf-b2409b1aa6f4
...
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: 9fa480e0-4967-4542-9390-d343dc5d04ae
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [CHG] Device 74:B5:87:92:0D:35 UUIDs: d0611e78-bbb4-4591-a5f8-487910ae4366
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [NEW] Primary Service (Handle 0xb64c)
        /org/bluez/hci0/dev_74_B5_87_92_0D_35/service0006
        00001801-0000-1000-8000-00805f9b34fb
        Generic Attribute Profile
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [NEW] Characteristic (Handle 0x006c)
        /org/bluez/hci0/dev_74_B5_87_92_0D_35/service0006/char0007
        00002a05-0000-1000-8000-00805f9b34fb
        Service Changed
...
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): [NEW] Descriptor (Handle 0xe4fc)
        /org/bluez/hci0/dev_74_B5_87_92_0D_35/service002d/char0036/desc0038
        00002900-0000-1000-8000-00805f9b34fb
        Characteristic Extended Properties
[agent] Authorize service 0000110d-0000-1000-8000-00805f9b34fb (yes/no): yes
Authorize service
[agent] Authorize service 0000110e-0000-1000-8000-00805f9b34fb (yes/no): yes
```

## OnOff Shim

A documenter