## Cr3dOv3r-攻击邮件

[github](https://github.com/D4Vinci/Cr3dOv3r)

- 检查目标邮件是否存在泄露，然后使用泄露的密码与网站进行核对。
- 检查您找到的目标凭据是否在其他网站/服务上重复使用。
- 检查您从 target/leaks 获得的旧密码是否仍在任何网站中使用。


## Usage

    usage: Cr3d0v3r.py [-h] [-p] [-np] [-q] email

    positional arguments:
      email       Email/username to check

    optional arguments:
      -h, --help  show this help message and exit
      -p          Don't check for leaks or plain text passwords.
      -np         Don't check for plain text passwords.
      -q          Quiet mode (no banner).
