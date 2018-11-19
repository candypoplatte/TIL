---
title: Content-Disposition 헤더
date: 2018-11-15 23:49:16
categories:
  - development
---

## Content-Disposition 헤더

HTTP Response 헤더의 종류. 브라우저에 Content가 `inline`으로 표시될지 `attachment`로 표시될지 여부를 나타낸다.

- `inline` : Web Page로, 혹은 Web Page 내에서 표시 (기본값)
- `attachment` : 로컬에 다운로드 & 저장 (대부분의 브라우저에서는 바로 다운로드가 되거나, "Save As" 다이얼로그가 표시됨)
  - `filename` : 파일 이름을 지정할 수 있음

```text
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"
```

## AWS S3

AWS S3에 저장된 File Object의 Metadata에 (`Content-Disposition`: `attachment`)가 포함되어 있으면, Object 다운로드 링크에 GET Request 시 Response에 `Content-Disposition` 헤더가 포함된다.

- 파일 업로드 시, `Content-Disposition` 헤더를 포함하여 업로드하면 됨
- 이미 저장된 파일일 경우, AWS Console 혹은 API를 활용하여 Metadata를 수정할 수 있음

## Django

`django-storages` 라이브러리를 사용하는 경우, `settings`에 `AWS_HEADERS` 혹은 `AWS_S3_OBJECT_PARAMETERS` 변수에 업로드 시 포함할 헤더를 지정할 수 있음

```python
# boto only
AWS_HEADERS = {
    'Content-Disposition': 'attachment'
}

# boto3 only
AWS_S3_OBJECT_PARAMETERS = {
    'Content-Disposition': 'attachment'
}
```

## Links

- [Content-Disposition 헤더](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition)
- [django-storages](https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html)