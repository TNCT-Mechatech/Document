# GPIO��p��������

- ## GPIO �Ƃ�

GPIO�́AGeneral Purpose Input/Output �̗��ł���BRaspberry Pi���܂ރR���s���[�^�ɊeGPIO�[�q�ɂ͓���̋@�\��������Ă���A���[�U�[�̈ӎv�Ŏ��R�ɓd�C�M���̓��o�͂𑀍�ł���B

# �{��

## pigpio

�ǂݕ���*�p�C�E�W�[�s�[�A�C�I�[*�ł���B������*�҂��҂�*�Ȃ�ĂւȂ��傱�Ȗ��O�ł͂Ȃ��B

- ### �T�v

pigpio �Ƃ� Raspberry Pi ��GPIO��p��������ɗp���郉�C�u�����ł���B���ׂ�����ł́AC++, Python�ɑΉ����Ă���B

- ### ����

������[pigpio�����T�C�g](https://abyz.me.uk/rpi/pigpio/)���Q�l�ɍs���B

1. **�{�̂̃C���X�g�[��**

�^�[�~�i���ňȉ��̃R�}���h�����s����B
```
wget https://github.com/joan2937/pigpio/archive/master.zip #github����pigpio�{�̂��_�E�����[�h
unzip master.zip   #�_�E�����[�h���� pigpio�{�̂���
cd pigpio-master   #�V�����f�B���N�g���Ɉړ�����B(�Ȃ��ꍇ�͍쐬���邱�ƁB->mkdir)
make
sudo make install  #�C���X�g�[��
```

����� pigpio �� Raspberry Pi �ɃC���X�g�[�������B

(Python �Ő�����������Ƃ��͂܂��ʂɃC���X�g�[�����K�v�Ȃ��̂�����Ƃ��Ȃ��Ƃ��B)

2. **���ϐ��̌p���ƒǉ�(ros2���g���ꍇ�̂�)**

���ɁAsudo���[�U�[��ros2 humble����F���ł���悤�Ɋ��ϐ���ݒ肷��K�v������B

�^�[�~�i���ňȉ��̃R�}���h�����s����B
```
nano /stc/environment
```

����ƁA�ȉ��̂悤�ȉ�ʂɂȂ�͂��ł���B
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

�����̓V�X�e���S�̂̃f�t�H���g�̊��ϐ����`����ꏊ�ł���B�����ݒ�ł� PATH ���ϐ��݂̂��L����Ă���B

��������������������B

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
LD_LIBRARY_PATH="/opt/ros/humble/lib:/usr/local/lib:/usr/lib"
PYTHONPATH="/opt/ros/humble/lib/python3.10/site-packages:/opt/ros/humble/local/lib/python3.10/dist-packages"
```

�������邱�Ƃɂ���āAroot�܂߁A���ׂẴ��[�U�[���A����������`LD_LIBRARY_PATH`��`PYTHONPATH`���g�����Ƃ��ł���悤�ɂȂ�B

�Ō�ɁAroot���[�U�[�Ƃ��Ẵm�[�h�̎��s�ɕK�v�ȋ@�\���g�����߂Ɋ��ϐ���ǉ����Ă����B

�^�[�~�i���ňȉ��̃R�}���h�����s����B

```
sudo visudo
```

����Ƃ��̂悤�ȉ�ʂɂȂ�B

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty

# This preserves proxy settings from user environments of root
# equivalent users (group sudo)
#Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"

# This allows running arbitrary commands, but so does ALL, and it means
# different sudoers have their choice of editor respected.
#Defaults:%sudo env_keep += "EDITOR"

#����...
```

������sudo�R�}���h�̓���⌠�����Ǘ�����d�v�Ȑݒ�t�@�C���ł���A���[�U�[��O���[�v��sudo���g���ăR�}���h�����s����ۂ̃��[�����L�q����Ă���B

�������������������B

```
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty
Defaults        env_keep += "PYTHONPATH"
Defaults        env_keep += "LD_LIBRARY_PATH"
Defaults        env_keep += "PATH"
Defaults        env_keep += "AMENT_PREFIX_PATH"
```

�ǉ����镔����`Defaults env_pty`�̉��̎l�݂̂ł���A���̂ق���`#`�R�����g�A�E�g���͏��������Ȃ��Ă悢�B

�ȏ�œ����͏I���ł���B���Ԃ�B