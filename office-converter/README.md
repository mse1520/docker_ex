# Office Converter
여러 종류의 문서 파일을 다른 형태의 문서로 변환해주는 api 서버입니다.
- hwp, hwpx 문서는 완벽히 작동하지 않습니다.

## 주요 기능 설정
- `libreoffice`를 이용해서 기본 문서를 변환합니다.
  ```sh
  # libreoffice를 이용한 파일 변경 명령어.
  soffice --headless --convert-to pdf ./input/test.pptx --outdir output
  ```
- `libreoffice-h2orestart`를 이용해서 hwp 문서 변환도 확장시킵니다.
- `fonts-nanum`, `fonts-noto-cjk` 등의 한글 폰트를 설치해서 한글문서의 텍스트 깨짐을 방지합니다.

## 부가 기능 설정
- `libreoffice-h2orestart`가 저장소에 존재하지 않을 경우 컨테이너 서버(Ubuntu) 버전 확인하기.