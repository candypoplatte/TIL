---
title: Django RunPython 실행할 때 Model을 직접 Import 하지 말자
date: 2018-11-12 23:49:16
categories:
  - development
---

`migrate` 커맨드로 Migration을 진행할 때, Migration 전/후로 실행할 코드가 있으면 함수를 작성하여 `operations` List에 추가한다.

**이 때 해당 함수가 Model을 사용한다면 Model Class를 직접 Import 하지 않는다.**

만약 해당 Model이 수정되었다면 문제가 된다. Migration 이전에 실행되어야하는 RunPython 코드가 새 버전의 Model 코드를 바라보게 되기 때문이다.

App Registry에는 이미 로딩된 설정이 올라가있을테니, App Registry에서 Model을 불러오면 된다!

```python
# NO
from test_app.models import TestModel

def forwards_func(apps, schema_editor):
    TestModel.objects.filter(field=True).update(field=False)

# YES
def forwards_func(apps, schema_editor):
    TestModel = apps.get_model("test_app", "TestModel")
    db_alias = schema_editor.connection.alias
    TestModel.objects.using(db_alias).filter(field=True).update(field=False)
```

## Links

- https://docs.djangoproject.com/en/2.1/ref/migration-operations/#runpython