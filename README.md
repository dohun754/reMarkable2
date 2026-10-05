# Installing Korean Font in reMarkable2 Paper Tablet

## Disclaimer
- This works in reMarkable2 firmware ver. 3.29.0.149 (as of Oct 6th)

## Step-by-Step Guide

### 1. Access reMarkable2 Tablet

reMarkable2와 PC를 USB 케이블을 통해 연결

SSH 클라이언트 (UNIX 내장 터미널은 기본 탑재) 통해 reMarkable2로 접속
- 참고: https://support.remarkable.com/s/article/Help
- `Settings > Help > Copyrights and licenses`에서 `root` 계정의 비밀번호와 IP를 찾을 수 있음

### 2. Download NotoKR Font

reMarkable2는 이미 Noto Sans의 TTF 포맷 폰트를 사용하고 있음.
동일 패밀리의 한글지원 버전 "Noto Sans KR"를 사용함. ([Noto Sans KR](https://fonts.google.com/noto/specimen/Noto+Sans+KR))

링크를 통해 압축 파일을 다운로드한 후 풀면 최상단에 `NotoSansKR-VariableFont_wght.ttf` 파일이 존재함.
이 파일은 여러가지 스타일이 한 파일 내에 들어있는 것인데 단일 스타일 파일을 모두 입력하는 것보다 용량을 덜 차지함 (용량 문제는 후술).

### 3. Now onto the Terminal!

reMarkable2은 그 자체로 리눅스 소프트웨어를 탑재하고 있으며, 파티션을 따로 관리할 수 있음.
단, 메인 파티션이 이미 97% 찬 상태로 배포됨.

따라서 폰트 디렉토리 자체를 `/home`으로 옮기고, Symbolic Link로 대체하여 용량 압박에서 벗어나야 함.

```bash
mv /usr/share/fonts/ttf ~/ttf
ln -sf /home/root/ttf /usr/share/fonts/ttf
```

### 4. Send Font File to reMarkable2

`scp`를 이용해 폰트 파일을 `~/ttf`로 전송한 뒤, 파일 권한을 설정함.

```bash
cd
chmod 644 ttf/NotoSansKR-VariableFont_wght.ttf
```

### 5. Font Application

단순히 파일만 넣는다고 해서 reMarkable2 시스템이 폰트를 읽어올 수는 없음. 아래 명령어를 통해 시스템이 폰트 파일을 읽어올 수 있도록 함.

```bash
fc-cache -fv
```

참고: `-f`는 필요는 없으나 공식 문서에서는 권장하고 있으며, `-v` 역시 불필요하나 터미널 상 어떤 디렉토리가 검색되는지 눈으로 보기 위함임.

```bash
fc-list | grep KR
```

아래와 같이 나오면 정상적으로 인식이 된 것임.

```bash
root@reMarkable:~/ttf# fc-list | grep KR
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=ExtraBold
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Light
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=SemiBold
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=ExtraLight
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Medium
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Regular
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Thin,Regular
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Bold
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin
/usr/share/fonts/ttf/NotoSansKR-VariableFont_wght.ttf: Noto Sans KR,Noto Sans KR Thin:style=Black
```

### 6. Reboot

```bash
systemctl restart xochitl
```
