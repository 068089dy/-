### 序列化
将model序列化：
比如，在Article表中，有'id', 'title', 'description', 'classification', 'tags', 'date', 'volume'这些字段，其中classification和tags是外键，
所以需要将其对应的序列化表示出来。
```
from rest_framework import serializers
from .models import Article, Classification, Tag


class ClassificationSerializer(serializers.ModelSerializer):
    class Meta:
        model = Classification
        fields = ('id', 'name', 'description', 'date')


class TagSerializer(serializers.ModelSerializer):
    class Meta:
        model = Tag
        fields = ('id', 'name', 'description', 'date')


class ArticleSerializer(serializers.ModelSerializer):
    classification = ClassificationSerializer(read_only=True)
    tags = TagSerializer(read_only=True)

    class Meta:
        model = Article
        fields = ('id', 'title', 'description', 'classification', 'tags', 'date', 'volume')

```
### 1.ViewSet
drf的ViewSet继承了
Viewset一般会和router一起使用，以达到一个标准的rest规范的效果，比如：
views.py
```
class ArticleViewSet(viewsets.ModelViewSet):
    """
    查看、编辑article
    """
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer
```
urls.py
```
from .views import ArticleListView, ArticleViewSet
from rest_framework import routers

router = routers.DefaultRouter()
router.register(r'article', ArticleViewSet)	#连接uri /users/和view userviewset


urlpatterns = [
    path('', include(router.urls)),
```
这样，访问/article/就可以获取到所有的article的json数据。/article/接口是一个标准的rest api，可以put，post，get。/article/id可以访问指定id的article
项。
### 2.APIView
drf的APIView继承了django的View
```

```
### Token 认证
1。添加app
```
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```
2。创建用户时生成token
```
# views.py
from rest_framework.authtoken.models import Token
from django.db.models.signals import post_save
from django.dispatch import receiver


# 创建user时生成Token
@receiver(post_save, sender=django.contrib.auth.models.User)
def create_auth_token(sender, instance=None, created=False, **kwargs):
    if created:
        Token.objects.create(user=instance)
```
3.如何获取token
法1：
使用rest框架自带的view

```
from rest_framework.authtoken import views

urlpatterns = [
    ...
    path('api-token-auth/', views.obtain_auth_token)
]
```
法2：
```
# views.py
from rest_framework.authtoken.views import ObtainAuthToken
from rest_framework.authtoken.models import Token
from rest_framework.response import Response


class CustomAuthToken(ObtainAuthToken):
    authentication_classes = ()

    def post(self, request, *args, **kwargs):
        serializer = self.serializer_class(data=request.data,
                                           context={'request': request})
        serializer.is_valid(raise_exception=True)
        user = serializer.validated_data['user']
        token, created = Token.objects.get_or_create(user=user)
        return Response({
            'token': token.key,
            'user_id': user.pk,
            'email': user.email
        })
        
# urls.py
path('api-token-auth/', views.CustomAuthToken.as_view())
```
这样，在post参数中添加username和password即可获取到token，header中要加Content-Type ： application/x-www-form-urlencoded。
4.使用token请求
``` python
# 在通用视图中添加token认证
from rest_framework.authentication import TokenAuthentication


class ArticleListView(
    generics.ListAPIView,
    mixins.CreateModelMixin
):
    authentication_classes = (TokenAuthentication, )
    ...
    # post请求认证
    # @wrap_permission(permissions.IsAdminUser)
    def post(self, request, *args, **kwargs):
        self.serializer_class = FactorySerializer.get_serializer(Article, attr_exclude=('volume', ))
        return self.create(request, *args, **kwargs)
```
客户端请求时，要在请求头header中加入：
```
Authorization ： Token 9c92543f96b36ac6de64a48e06b1a65ada83a38e
```
### 通用视图中为不同方法设置不同的认证
写一个装饰器
``` python
from functools import update_wrapper


def wrap_permission(*permissions, validate_permission=True):
    """custom permissions for special route"""
    def decorator(func):
        def wrapper(self, request, *args, **kwargs):
            self.permission_classes = permissions
            if validate_permission:
                self.check_permissions(request)
            return func(self, request, *args, **kwargs)
        return update_wrapper(wrapper, func)
    return decorator
```
假如要为post请求设置权限
```
...
    @wrap_permission(permissions.IsAdminUser)
    def post(self, request, *args, **kwargs):
        self.serializer_class = FactorySerializer.get_serializer(Article, attr_exclude=('volume', ))
        return self.create(request, *args, **kwargs)
...
```
