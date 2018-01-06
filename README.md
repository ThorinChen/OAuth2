### 基本说明
基于SpringSecurity和OAuth2结合,架构出的安全验证中心,

_ _ _

### grant_type
说明OAuth2支持的grant_type(授权方式)与功能
`authorization_code` -- 授权码模式(即先登录获取code,再获取token)
`password` -- 密码模式(将用户名,密码传过去,直接获取token)
`refresh_token` -- 刷新access_token
`implicit` -- 简化模式(在redirect_uri 的Hash传递token; Auth客户端运行在浏览器中,如JS,Flash)
`client_credentials` -- 客户端模式(无用户,用户向客户端注册,然后客户端以自己的名义向'服务端'获取资源)


_ _ _

### 运行后
尝试不使用任何验证信息来直拉访问资源：http://localhost:8080/SpringSecurityOAuth2/user/，将得到401。
现在我们获取头。选择HTTP方法为 POST，
Authorization Type：Basic Auth ，（用户名和密码为鉴权服务器配置的客户端ID和serct）
URL：http://localhost:8080/SpringSecurityOAuth2/oauth/token?grant_type=password&username=bill&password=abc123 ，
然后再将客户端凭据 [my-trusted-client/secret]添加到授权头。
点击"update request"(更新请求)，发送POST请求后，您会在响应中收到访问令牌(access-token)，以及刷新令牌(refresh-token)。

保存这些令牌在需要它们时。现在可以使用这个访问令牌[有效期为2分钟]来访问资源。现在我们再使用这个 token 来访问资源，
把它添加到URL中如：http://localhost:8080/SpringSecurityOAuth2/user/?access_token=7fbb77ae-3d8f-4d78-b8de-3222353f680b 

2分钟后，访问令牌被过期，那么进一步的资源请求将失败。

我们需要一个新的访问令牌。触发一个 post 以后用刷新令牌来获得一个新的访问令牌。
  请求URL：http://localhost:8080/SpringSecurityOAuth2/oauth/token?grant_type=refresh_token&refresh_token=fefcf12c-2683-4f1a-a446-941666dcfe23
  
使用这个新的访问令牌(c8edfa2f-d2aa-4f1b-81e1-32df3fefe9a8)继续访问资源。把它添加到URL中如：http://localhost:8080/SpringSecurityOAuth2/user/?access_token=be5c7dec-ae17-403d-ab66-86cf5262f159 得到结果

新令牌(Refresh-token)也会过期[10分钟]。在这之后，您会看到刷新请求失败。

这意味着您需要刷新申请新的访问令牌，如第2步中。


