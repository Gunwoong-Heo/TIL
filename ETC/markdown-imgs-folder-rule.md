# 마크다운file 이미지 폴더 naming 규칙

### 기본 규칙
* IDE 사이드바 에서는 최대한 md파일만 보여야한다.
* 이미지폴더는 마크다운파일이름/자유형식의이미지이름.jpg 이어야 한다.
* 파일을+이미지폴더 drag&drop 형식으로 쉽게 옮기기가 가능해야한다.
* IDE 탐색기에서 oneClick으로 파일끼리 쉽게 이동하면서 볼수 있어야 한다.
* md파일이 폴더를 넘나들며 이동하여도 md파일 본문의 이미지 경로를 수정하지 않아야한다.
* markdown-tistory 로 write&update 가 되어야 한다.

<br/>

### md파일 구조 예시
* 카테고리/md파일이름.md
  * tistory/Spring/Test/assertThrows.md  

<br/>

### img파일 구조 예시
* 카테고리/imgs/md파일이름/숫자(img파일이름).확장자
  * tistory/Spring/Test/imgs/assertThrows/01(error_sample).jpg
  * tistory/Spring/Test/imgs/assertThrows/01A(error_sample).jpg    <-추후 추가시 `알파벳` 으로 구분
  * tistory/Spring/Test/imgs/assertThrows/02(error_sample).jpg

<br/>

### VSCode extension 설정 (paste image)
* Paste Image: Path : `${currentFileDir}/imgs/${currentFileNameWithoutExt}`