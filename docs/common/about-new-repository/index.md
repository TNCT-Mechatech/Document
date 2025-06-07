


# ROS2 C++ �v���W�F�N�g �`�[���J���^�p�}�j���A��

---

## �ڎ�

1. [���|�W�g���쐬�E������](#���|�W�g���쐬������)
2. [Raspberry Pi ���ł̃Z�b�g�A�b�v](#raspberry-pi-���ł̃Z�b�g�A�b�v)
3. [SSH���̐�����GitHub�o�^](#ssh���̐�����github�o�^)
4. [�����[�g���|�W�g���̃N���[��](#�����[�g���|�W�g���̃N���[��)
5. [���|�W�g�������R�~�b�g](#���|�W�g�������R�~�b�g)
6. [.gitignore�̐ݒ��](#gitignore�̐ݒ��)
7. [�R�~�b�g�Epull�Epush�̊�{�菇](#�R�~�b�gpullpush�̊�{�菇)
8. [�u�����`�^�p��Pull Request](#�u�����`�^�p��pull-request)
9. [�g���u���V���[�e�B���O](#�g���u���V���[�e�B���O)
10. [�^�p���[���܂Ƃ�](#�^�p���[���܂Ƃ�)

---

## ���|�W�g���쐬�E������

### 1.1 GitHub�Ń��|�W�g����V�K�쐬

1. [GitHub](https://github.com/)�Ƀ��O�C�����A�uNew repository�v���N���b�N�B
2. Repository name ����́i��: `robot-control`�j
3. �uInitialize this repository with a README�v��**�`�F�b�N���O��**�i�󃊃|�W�g�������j
4. Public / Private ��I��
5. �쐬��A���|�W�g����SSH�܂���HTTPS URL���T����B

---

## Raspberry Pi ���ł̃Z�b�g�A�b�v

### 2.1 Git�̃C���X�g�[���i����̂݁j

```bash
sudo apt update
sudo apt install git
````

### 2.2 ���[�U�[���̐ݒ�i����̂݁j

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

---

## SSH���̐�����GitHub�o�^

### 3.1 �V����SSH�����쐬

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# ���̂܂�Enter�A�ł�OK�i�p�X�t���[�Y�͔C�Ӂj
```

### 3.2 ���J���̊m�F��GitHub�o�^

```bash
cat ~/.ssh/id_ed25519.pub
# �o�͂��ꂽ1�s�S�����R�s�[
```

* GitHub �� �E��A�C�R�� �� Settings �� SSH and GPG keys �� New SSH key
* Title�͎��R�AKey���ɃR�s�[�������J����\��t���A�uAdd SSH key�v

---

## �����[�g���|�W�g���̃N���[��

### 4.1 �v���W�F�N�g�̕ۑ���f�B���N�g���Ɉړ�

```bash
cd ~/your_workspace/
```

### 4.2 �N���[��

```bash
git clone git@github.com:YourUserName/robot-control.git
cd robot-control
```

---

## ���|�W�g�������R�~�b�g

### 5.1 �v���W�F�N�g�t�@�C���̔z�u

�i��F`include`, `src`, `CMakeLists.txt`�Ȃǂ����̃f�B���N�g�������ɔz�u�j

### 5.2 .gitignore�̍쐬

```bash
nano .gitignore
# �K�v�ȓ��e���L�ځi��͉��L�j
```

### 5.3 �ŏ��̃R�~�b�g��push

```bash
git add .
git commit -m "����R�~�b�g: �v���W�F�N�g�����\��"
git branch -M main
git push -u origin main
```

---

## .gitignore�̐ݒ��

```gitignore
/build/
/install/
/log/
*.swp
*.swo
*.pyc
.vscode/
.DS_Store
```

---

## �R�~�b�g�Epull�Epush�̊�{�菇

### 7.1 ��ƑO��pull�i���l�̍X�V���擾�j

```bash
git pull origin main
```

### 7.2 �t�@�C���̕ҏW

�iVSCode�Ń����[�g�ڑ����ҏW�����j

### 7.3 �ύX�̊m�F

```bash
git status
```

### 7.4 �ύX�̒ǉ�

```bash
git add .
# �܂���
git add �t�@�C����
```

### 7.5 �R�~�b�g

```bash
git commit -m "�ύX���e���Ȍ��ɋL�q"
```

### 7.6 push

```bash
git push origin �u�����`��
# ��Fgit push origin main
```

---

## �u�����`�^�p��Pull Request

### 8.1 �V�K�u�����`�̍쐬

```bash
git checkout -b feature/xxxx
```

### 8.2 �ύX���e��push

```bash
git push origin feature/xxxx
```

### 8.3 Pull Request�̍쐬

1. GitHub�Ń��|�W�g���y�[�W���J��
2. `feature/xxxx`�u�����`�ŁuCompare & pull request�v���N���b�N
3. ���r���[���󂯁A���F���ꂽ��main�Ƀ}�[�W

### 8.4 main�u�����`�̍X�V��pull

```bash
git checkout main
git pull origin main
```

---

## �g���u���V���[�e�B���O

### SSH�́uPermission denied (publickey)�v�G���[���o���ꍇ

* �����쐬�������āA\*\*���[�U�[�́uSSH and GPG keys�v\*\*�ɍēo�^���邱��
* Deploy key�i�ǂݎ���p�j�ɓo�^���Ȃ�����

### �uproject�v�f�B���N�g������push���Ă��܂����ꍇ

* �K�v�Ȃ�GitHub�̃��|�W�g������x�폜���A�f�B���N�g��������`include`��`src`������`�ō�push����

### SSH����ڑ����́uauthenticity of host�v���b�Z�[�W

* �t�B���K�[�v�����g��GitHub�����ƈ�v���Ă����`yes`��OK

---

## �^�p���[���܂Ƃ�

* **�R�~�b�g�Epush/pull�͕K��Raspberry Pi��œ��ꂵ�Ď��s**
* **main�u�����`�ɒ���push�֎~�A�K��feature�u�����`��Pull Request�ŉ^�p**
* **��ƑO�͕K��pull���čŐV��**
* **�R���t���N�g���o���ꍇ�͗���������`git status`/`git diff`�œ��e���m�F���A�C�����邱��**
* **.gitignore�����A�r���h��������s�v�t�@�C���̓��|�W�g���Ɋ܂߂Ȃ�**
* **README.md���ɃZ�b�g�A�b�v�菇�A���[���A�g���u������𐏎��ǋL����**

---

## �悭�g��Git�R�}���h�܂Ƃ�

| �R�}���h                        | �Ӗ�                      |
| --------------------------- | ----------------------- |
| git pull origin main        | �����[�gmain���擾���Ď茳��main�ɔ��f |
| git checkout -b feature/xxx | �V�����u�����`������Ă��̂܂ܐ؂�ւ�     |
| git add .                   | �ύX�����S�Ẵt�@�C����ǉ�          |
| git commit -m "�R�����g"        | �ǉ������R�~�b�g�i�R�����g�͕K������j    |
| git push origin feature/xxx | ���̃u�����`�������[�g�փA�b�v���[�h      |
| git checkout main           | main�u�����`�ɖ߂�             |
| git branch                  | �����݂���u�����`�ꗗ��\��          |
| git status                  | ���̕ύX�󋵂��m�F               |

