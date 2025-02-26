# Dial Plan Recording

## Setup Recording Directory

Ensure that the recording directory exists:

```sh
mkdir -p /var/spool/asterisk/monitor/
chmod -R 777 /var/spool/asterisk/monitor/
# OR
chown -R asterisk:asterisk /var/spool/asterisk/monitor/
```

Verify the directory permissions:

```sh
ls -lh /var/spool/asterisk/monitor/
```

## Configure `extensions.conf`

Edit your `extensions.conf` file and select your context. Then, add the following dial plan to enable call recording:

```plaintext
exten => _X.,1,Set(CALLFILENAME=${CALLERID(num)}-${EXTEN}-${STRFTIME(${EPOCH},,%Y-%m-%d-%H-%M-%S)})
exten => _X.,n,MixMonitor(/var/spool/asterisk/monitor/${CALLFILENAME}.wav)
exten => _X.,n,Dial(PJSIP/${EXTEN},30)
```

## Testing the Configuration

After applying the configuration, reload Asterisk and test by making a call:

```sh
asterisk -rvvv
```

Make a test call and check if the recording is saved in `/var/spool/asterisk/monitor/`.


![image](https://github.com/user-attachments/assets/05e8e387-6738-48a0-81a4-14c07ffc6468)

![image](https://github.com/user-attachments/assets/c09b9fb6-db81-43ca-865c-106671457bb0)

