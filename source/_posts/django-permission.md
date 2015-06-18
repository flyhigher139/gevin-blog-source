---
layout: post
title: Django权限机制的实现
date: 2015-06-17 16:42:01
category: tech
tags:
  - django
  - python
description: 权限机制能够约束用户行为，控制页面的显示内容，也能使API更加安全和灵活；用好权限机制，能让系统更加强大和健壮。因此，基于Django的开发，理清Django权限机制是非常必要的
---

![django-permission](http://7d9q7a.com1.z0.glb.clouddn.com/blog4041-34f7e1442009992f.jpg)



>本文涉及的技术，已应用于我基于django 1.8+ 开发的博客系统——[MayBlog](https://github.com/flyhigher139/mayblog)，欢迎交流。

##1. Django权限机制概述

权限机制能够约束用户行为，控制页面的显示内容，也能使API更加安全和灵活；用好权限机制，能让系统更加强大和健壮。因此，基于Django的开发，理清Django权限机制是非常必要的。

###1.1 Django的权限控制

Django用user, group和permission完成了权限机制，这个权限机制是将属于model的某个permission赋予user或group，可以理解为全局的权限，即如果用户A对数据模型（model）B有可写权限，那么A能修改model B的所有实例（objects）。group的权限也是如此，如果为group C 赋予model B的可写权限，则隶属于group C 的所有用户，都可以修改model B的所有实例。

这种权限机制只能解决一些简单的应用需求，而大部分应用场景下，需要更细分的权限机制。以博客系统为例，博客系统的用户可分为『管理员』、『编辑』、『作者』和『读者』四个用户组；博客系统管理员和编辑具有查看、修改和删除所有的文章的权限，作者只能修改和删除自己写的文章，而读者则只有阅读权限。管理员、编辑和读者的权限，我们可以用全局权限做控制，而对于作者，全局权限无法满足需求，仅通过全局权限，要么允许作者编辑不属于自己的文章，要么让作者连自己的文章都无法修改。

上述的应用场景，Django自带的权限机制无法满足需求，需要引入另一种更细的权限机制：`对象权限（object permission）`。

Object Permission是一种对象颗粒度上的权限机制，它允许为每个具体对象授权。仍沿用最开始的例子，如果model B有三个实例 B1，B2 和B3，如果我们把B1的可写权限赋予用户A，则A可以修改B1对象，而对B2，B3无法修改。对group也一样，如果将B2的可写权限赋予group C，则隶属于group C的所有用户均可以修改B2，但无法修改B1和B3。结合Django自带权限机制和object permission，博客系统中作者的权限控制迎刃而解：系统全局上不允许作者编辑文章，而对于属于作者的具体文章，赋予编辑权限即可。

Django其实包含了object permission的框架，但没有具体实现，object permission的实现需要借助第三方app[ django-guardian](https://github.com/lukaszb/django-guardian)，我们在开发中用调用django guradian封装好的方法即可。

###1.2 Django的权限项

Django用permission对象存储权限项，每个model默认都有三个permission，即add model, change model和delete model。例如，定义一个名为『Car』model，定义好Car之后，会自动创建相应的三个permission：`add_car`, `change_car`和`delete_car`。Django还允许自定义permission，例如，我们可以为Car创建新的权限项：`drive_car`, `clean_car`, `fix_car`等等

需要注意的是，permission总是与model对应的，如果一个object不是model的实例，我们无法为它创建/分配权限。

##2. Django 自带权限机制的应用

###2.1 Permission

如上文所述，Django定义每个model后，默认都会添加该model的add, change和delete三个permission，自定义的permission可以在我们定义model时手动添加：

```python
class Task(models.Model):
    ...
    class Meta:
        permissions = (
            ("view_task", "Can see available tasks"),
            ("change_task_status", "Can change the status of tasks"),
            ("close_task", "Can remove a task by setting its status as closed"),
        )
```

每个permission都是`django.contrib.auth.Permission`类型的实例，该类型包含三个字段`name`, `codename` 和 `content_type`，其中 `content_type`反应了permission属于哪个model，`codename`如上面的`view_task`，代码逻辑中检查权限时要用， `name`是permission的描述，将permission打印到屏幕或页面时默认显示的就是`name`

在model中创建自定义权限，从系统开发的角度，可理解为创建系统的内置权限，如果需求中涉及到用户使用系统时创建自定义权限，则要通过下面方法：

```python
from myapp.models import BlogPost
from django.contrib.auth.models import Permission
from django.contrib.contenttypes.models import ContentType

content_type = ContentType.objects.get_for_model(BlogPost)
permission = Permission.objects.create(codename='can_publish',
                                       name='Can Publish Posts',
                                       content_type=content_type)
```


###2.2 User Permission管理

User对象的user_permission字段管理用户的权限：

```python
myuser.user_permissions = [permission_list]
myuser.user_permissions.add(permission, permission, ...) #增加权限
myuser.user_permissions.remove(permission, permission, ...) #删除权限
myuser.user_permissions.clear() #清空权限

##############################################################
# 注：上面的permission为django.contrib.auth.Permission类型的实例
##############################################################
```

检查用户权限用`has_perm()`方法：

```python
myuser.has_perm('myapp.fix_car')
```

`has_perm()`方法的参数，即permission的`codename`，但传递参数时需要加上model 所属app的前缀，格式为`<app label>.<permission codename>`。

无论permission赋予user还是group，`has_perm()`方法均适用

**附注：**

`user.get_all_permissions()`方法列出用户的所有权限，返回值是permission name的list
`user.get_group_permissions()`方法列出用户所属group的权限，返回值是permission name的list

###2.3 Group Permission管理

group permission管理逻辑与user permission管理一致，group中使用permissions字段做权限管理：

```python
group.permissions = [permission_list]
group.permissions.add(permission, permission, ...)
group.permissions.remove(permission, permission, ...)
group.permissions.clear()
```

权限检查：

依然使用`user.has_perm()`方法。



###2.4 `permission_required` 装饰器

权限能约束用户行为，当业务逻辑中涉及到权限检查时，decorator能够分离权限验证和核心的业务逻辑，使代码更简洁，逻辑更清晰。permission的decorator为`permission_required`：

```python
from django.contrib.auth.decorators import permission_required

@permission_required('car.drive_car')
def my_view(request):
    ...
```

###2.5 Template中的权限检查

Template中使用全局变量`perms`存储当前用户的所有权限，权限检查可以参考下面例子：

```
{% if perms.main.add_page %}
            <li class="dropdown">
              <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Pages <span class="caret"></span></a>
              <ul class="dropdown-menu" role="menu">
                <li><a href="{% url 'main:admin_pages' %}">All Pages</a></li>
                <li><a href="{% url 'main:admin_page' %}">New Page</a></li>
                <li><a href="{% url 'main:admin_pages' %}?draft=true">Drafts</a></li>
              </ul>
            </li>
{% endif %}
```

##3. 基于Django-guardian的object permission的应用

[Django-guardian](https://github.com/lukaszb/django-guardian)基于django的原生逻辑扩展了django的权限机制，应用django-guardian后，可以使用django-guardian提供的方法以及django的原生方法检查全局权限，django-guardian提供的object permission机制使django的权限机制更加完善。


django-guardian详细的使用文档请参考官方文档，其object permission常用方法如下：

```python
from guardian.shortcuts import assign_perm, get_perms
from guardian.core import ObjectPermissionChecker
from guardian.decorators import permission_required
```

###3.1 添加object permission

添加object permission使用`assign_perm()`方法，如为用户添加对mycar对象的drive_car权限：

```python
assign_perm('myapp.drive_car', request.user, mycar) 
```

`assign_perm()`方法也可用于group

```python
assign_perm('myapp.drive_car', mygroup, mycar) 
```

###3.2 权限检查

####3.2.1 Global permission

`get_perms()`方法用于检查用户的“全局权限”（global permission），与`user.has_perm()`异曲同工，如：

```python
#############################
# It works! 
#############################
 if not 'main.change_post' in get_perms(request.user, post):
     raise HttpResponse('Forbidden')

#############################
# It works, too!
#############################
if not request.user.has_perm('main.change_post')
    return HttpResponse('Forbidden')
```

例子中虽然把post object作为参数传给get_perms()方法，但它只检查user的全局权限中是否有`main.change_post`权限，很多情况下可用原生的`user.has_perm`取代，但user和group均可作为`get_perms()`的传入参数，某些情况下可以使代码更简洁。

####3.2.2 Object permission

Django-guardian中使用`ObjectPermissionChecker`检查用户的object permission，示例如下：

```python
checker = ObjectPermissionChecker(request.user)
print checker.has_perm('main.change_post', post)
```


###3.3 permission_required装饰器

`guardian.decorators.permission_required`是django-guardian权限检查的decorator，既可以检查全局权限，又可以检查对象权限（object permission），其中，`accept_global_perms`参数指出是否检查user的global permission，如：

```python
from guardian.decorators import permission_required

class DeletePost(View):
    @method_decorator(permission_required('main.delete_post', 
                            (models.Post, 'id', 'pk'), 
                            accept_global_perms=True))
    def get(self, request, pk):
        try:
            pk = int(pk)
            cur_post = models.Post.objects.get(pk=pk)
            is_draft = cur_post.is_draft

            url = reverse('main:admin_posts')
            if is_draft:
                url = '{0}?draft=true'.format(url)    
            cur_post.delete()
        except models.Post.DoesNotExist:
            raise Http404

        return redirect(url)
```

**注：**

decorator中的`(models.Post, 'id', 'pk')`部分，用于指定object实例，如果忽略这个参数，则不论accept_global_perms值为True还是False，均仅仅检查全局权限。

##4. 结论

Django原生提供了一种简单的全局权限（global permission）控制机制，但很多应用场景下，对象权限（object permission）更加有用；django-guardian是目前比较活跃的一个django extension，提供了一种有效的object permission控制机制，与django原生机制一脉相承，推荐使用。