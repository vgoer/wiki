<center>ORM对象关系映射</center>



[toc]





## ORM 对象关系映射

> 通过 Eloquent 提供了简洁且强大的数据库关联功能。







### 1. 一对一

> 1. 一对一关联（One-to-One Relationship）： 假设我们有两个模型：User 和 Phone。一个用户只拥有一个电话号码。

```php
# User 模型中定义关联方法：

public function phone(): HasOne
{
    return $this->hasOne(Phone::class);
}
```

> 使用

```php
// 获取用户的电话号码
$user = User::find(1);
$phone = $user->phone;
```

```php
// 获取用户电话  findOrFail查id并过滤
$user = User::query()->findOrFail(1,['id','name']);
$profile = $user->profile;
```



> 反向关联

```php
# Phone 模型中定义反向关联方法：
public function user()
{
    return $this->belongsTo(User::class);
}
```





### 2. 一对多

> 1. 一对多关联（One-to-Many Relationship）： 假设我们有两个模型：Post 和 Comment。一篇文章可以有多个评论。

```php
# Post 模型中定义关联方法：
public function comments()
{
    return $this->hasMany(Comment::class);
}

// 一对多 指定别名 article_id
public function posts() : HasMany
{
    return $this->hasMany(Post::class,'article_id');
}
```

```php
# Comment 模型中定义反向关联方法：
public function post()
{
    return $this->belongsTo(Post::class);
}
```

> 使用

```php
// 获取一篇文章的评论
$post = Post::find(1);
$comments = $post->comments;
// 获取用户的所有文章
$user = User::find(1);
$posts = $user->posts;

$user = User::query()->findOrFail(1);

$posts = $user->posts;
dd($posts); //查出这个用户的所有记录
```





### 3. 多对多

> 1. 多对多关联（Many-to-Many Relationship）： 假设我们有两个模型：User 和 Role。一个用户可以属于多个角色，一个角色也可以被多个用户拥有。

```php
#  User 模型中定义关联方法：
public function roles()
{
    return $this->belongsToMany(Role::class);
}

```

```php
# Role 模型中定义反向关联方法：
public function users()
{
    return $this->belongsToMany(User::class);
}
```

> 使用

```php
// 获取一个角色的所有用户
$role = Role::find(1);
$users = $role->users;
```





### 4. 多态关联

> 1. 多态关联（Polymorphic Relationship）： 假设我们有两个模型：Post 和 Video。评论可以属于这两个模型中的任何一个。

```php
// Post 模型
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}

// Video 模型
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}
```

```php
# Comment 模型中定义反向关联方法：
public function commentable()
{
    return $this->morphTo();
}

```

> 使用

```php
1. 创建

创建 posts 表用于存储文章信息，包括 id、title 等字段。
创建 videos 表用于存储视频信息，包括 id、title 等字段。
创建 comments 表用于存储评论信息，包括 id、content 等字段，还需要创建 commentable_id 和 commentable_type 用于多态关联。
    
2. 模型
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}

class Video extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}

class Comment extends Model
{
    public function commentable()
    {
        return $this->morphTo();
    }
}

3. 使用
// 创建一篇文章及其评论
$post = Post::create(['title' => 'Hello, World!']);
$comment1 = $post->comments()->create(['content' => 'Great post!']);
$comment2 = $post->comments()->create(['content' => 'Nice article!']);

// 创建一个视频及其评论
$video = Video::create(['title' => 'Introduction to Laravel']);
$comment3 = $video->comments()->create(['content' => 'Informative video']);
$comment4 = $video->comments()->create(['content' => 'Well-explained tutorial']);

// 查询一篇文章的所有评论
$post = Post::find(1);
$comments = $post->comments;

// 查询一个视频的所有评论
$video = Video::find(1);
$comments = $video->comments;
```



















