---
title: 링크드인에 자료 업로드 시 한글이 다 사라져 버릴 때
date: 2019-01-01
categories:
  - development
---

최근 링크드인에 발표 자료를 공유할 일이 있었다. 그런데 업로드만 하면 모든 글자가 사라져 버려서 안절부절하던 중 아래의 블로그 포스트를 보게 되었다. (감사합니다. item4)

[SlideShare에서 자국어 폰트 사용하기 - item4](https://item4.github.io/2016-10-31/Way-to-Use-Homeland-Fonts-on-SlideShare)

요약하자면, 아래의 커맨드를 입력 후 실행하면 된다. (이유는 위 블로그 참고. 사실 완벽하게 이해하지는 않음)

```bash
LANG=C LC_ALL=C sed -i '' s'|/Registry (Adobe) /Ordering (Korea1) /Supplement [0-9]|/Registry(Adobe) /Ordering(Identity) /Supplement 0|g' /path/to/pdf.pdf
```