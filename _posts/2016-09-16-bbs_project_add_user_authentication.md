---
layout: post
title: 【laravel分享】一步一步构建类PHPHub网站：加入用户认证
---

# 发帖之前要注册

作为一个论坛，并不是所有人都能够发贴，最起码需要登陆之后才能够发贴。所以初级的论坛系统就需要增加一个用户登陆后再发贴的功能。那么，第二个阶段的需求就出来了。

* 增加用户认证
* 增加注册、登陆
* 增加只有登陆才能够发贴
* 增加只能删除自己的Post或Comment

Laravel提供了非常完整的用户认证框架，可以直接去使用。具体可以参考[文档](https://laravel.com/docs/master/authentication)

测试代码如下：

```
/**
 * @test
 */
public function 注册()
{
    $userData = [
        'name'      =>  'wang',
        'email'     =>  'wang@wang.com',
        'password'  =>  '123456',
        'password_confirmation' =>  '123456',//容易忘记写这个
    ];
    $this->visit('/register')
        ->submitForm('Register',$userData)
        ->seeCredentials($userData)
        ->seePageIs('/')
        ->seeIsAuthenticated();
}
/**
 * @test
 */
public function 登陆()
{
    $userData = [
        'email'     =>  'wang@wang.com',
        'password'  =>  '123456',
    ];
    $user = factory(User::class)->create([
        'email' => $userData['email'],
        'password'  => bcrypt($userData['password']),
    ]);
    $this->visit('/login')
        ->submitForm('Login',$userData)
        ->seeCredentials($userData)
        ->seePageIs('/')
        ->seeIsAuthenticated();
}
/**
 * 由于前台logout采用js提交，暂时就不理了
 */
public function 退出登陆()
{
    $userData = [
        'email'     =>  'wang@wang.com',
        'password'  =>  '123456',
    ];
    $user = factory(User::class)->create([
        'email' => $userData['email'],
        'password'  => bcrypt($userData['password']),
    ]);
    $this->visit('/login')
        ->submitForm('Login',$userData)
        ->seePageIs('/')
        ->seeIsAuthenticated();
    $this->visit('/')
        ->submitForm('logout-form');
    $this->dontSeeIsAuthenticated();
}
```

# 在测试中模拟登陆

Laravel的测试提供了一个actingAs(User)的方法。通过这个方法，可以方便的模拟用户登陆。在需要用户登陆地方，使用这个方法，就可以对用户进行认证了。

```
/**
 * @test
 */
public function 新建Post()
{
    $data = [
        'title'     =>  'this is title',
        'content'   =>  'blog content',
    ];
    $this->actingAs($this->user)
        ->visit(route('post.create'))
        ->submitForm('create',$data);
    $this->see('create success');
    $this->seeInDatabase('posts',$data);
 }
```

# 代码地址

以上内容的代码，请[点击](https://github.com/polly3d/bbs/tree/2-second-step)前往。