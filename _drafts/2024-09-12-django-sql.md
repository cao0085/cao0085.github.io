---
title: "Django "
date: 2024-09-12
categories: setup-basics
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
#####

<!-- 正文 -->

先做核心 輸入網址->回應 
urls.py
```python
from django.contrib import admin
from django.urls import path
from . import views  # 引入 views.py

urlpatterns = [
    path('admin/', admin.site.urls),
    path('show_data/', views.show_data, name='show_data'),  # 添加 show_data 路徑
]
```
views.py
```python
from django.http import HttpResponse


def show_data(request):
    return HttpResponse("This is the show_data page.")
```

---

測試Django-> MySQL
檢查 mysqlclient==2.2.4 
setting.py
```python
    'mysql': {  # 添加 MySQL 資料庫配置
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.getenv('DJANGO_DB_NAME'),  # 使用環境變數獲取資料庫名稱
        'USER': os.getenv('DJANGO_DB_USER'),  # 使用環境變數獲取資料庫使用者
        'PASSWORD': os.getenv('DJANGO_DB_PASSWORD'), }
```

python manage.py migrate

views.py
```python
from django.http import HttpResponse
from django.db import connection

def show_data(request):
    # 使用 Django 的資料庫連接執行 SQL 查詢
    with connection.cursor() as cursor:
        cursor.execute("SHOW DATABASES;")
        databases = cursor.fetchall()

    # 將資料庫名稱列表轉換成字串
    databases_list = ", ".join([db[0] for db in databases])

    # 將結果顯示在頁面上
    return HttpResponse(f"Databases: {databases_list}")
```

change defultDB

```python
DATABASES = {
    'default': {  # 將默認資料庫設置為 MySQL
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.getenv('DJANGO_DB_NAME'),
        'USER': os.getenv('DJANGO_DB_USER'),
        'PASSWORD': os.getenv('DJANGO_DB_PASSWORD'),
        'HOST': os.getenv('DJANGO_DB_HOST'),
        'PORT': '3306',
    }
}
```



