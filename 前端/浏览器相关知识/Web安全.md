Q：前端常见的安全问题有哪些？

- XSS（Cross Site Scripting，跨站脚本攻击）
  - 危害：盗取用户cookie信息
  - 预防：
    - 对cookie进行强控制，对敏感的cookie增加http-only限制，让js获取不到cookie的内容
    - 对用户输入的内容进行验证和替换
```
& 替换为：&amp;
< 替换为：&lt;
> 替换为：&gt;
” 替换为：&quot;
‘ 替换为：&#x27;
/ 替换为：&#x2f;
```

- CSRF（Cross-site request forgery，跨站请求伪造）
  - 基础：浏览器会共享本地cookie
  - 预防：加入各个层级的权限验证，涉及交易需要输入密码或者指纹等
