### from django.shortcuts

```
from django.shortcut import render, redirect, get_object_or_404, get_list_or_404
```

* render()

* redirect()

* get_object_or_404() : 모델에서 get()을 호출 / 없는경우 DoesNotExist 예외 대신 Http 404를 raise

  ```
  article = get_object_or_404(Article, pk=pk)
  ```

* get_list_or_404()

### from django.views.decorators.http

request가 조건을 충족시키지 못하면 HttpResponseNotAllowed 반환(405 error)

```
from django.views.decorators.html import require_http_methods(), require_POST(), require_safe()
```

* require_http_methods() : 특정한 method요청에 대해서만 허용

  ```
  @require_http_methods(['GET','POST'])
  ```

* require_POST()

  ```
  @require_POST
  ```

* require_safe()

* require_GET() - 안씀(위에있는 safe 씀)