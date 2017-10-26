---
layout: post
title:  "SpringMVC-day02"
categories: SpringMVC
tags: SpringMVC
author: Jason
excerpt_separator: "###"
---

* 目录
{:toc}

## 高级参数绑定

### 绑定数组

```
/**
 * 包装类型 绑定数组类型，可以使用两种方式，pojo的属性接收，和直接接收
 * 
 * @param queryVo
 * @return
 */
@RequestMapping("queryItem")
public String queryItem(QueryVo queryVo, Integer[] ids) {

	System.out.println(queryVo.getItem().getId());
	System.out.println(queryVo.getItem().getName());

	System.out.println(queryVo.getIds().length);
	System.out.println(ids.length);

	return "success";
}
```

### 绑定List
1. List中存放对象，并将定义的List放在包装类QueryVo中

```
//用对象属性接受List集合
List<Items> itemList; 
```

2. JSP页面

```
<c:forEach items="${itemList }" var="item" varStatus="s">
<tr>
	<td><input type="checkbox" name="ids" value="${item.id}"/></td>
	<td>
		<input type="hidden" name="itemList[${s.index}].id" value="${item.id }"/>
		<input type="text" name="itemList[${s.index}].name" value="${item.name }"/>
	</td>
	<td><input type="text" name="itemList[${s.index}].price" value="${item.price }"/></td>
	<td><input type="text" name="itemList[${s.index}].createtime" value="<fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/>"/></td>
	<td><input type="text" name="itemList[${s.index}].detail" value="${item.detail }"/></td>
	
	<td><a href="${pageContext.request.contextPath }/itemEdit.action?id=${item.id}">修改</a></td>

</tr>
</c:forEach>
```

## RequestMapping

1. URL路径映射
> @RequestMapping(value="item")或@RequestMapping("/item"）
2. 添加在类上
> 在class上添加@RequestMapping(url)指定通用请求前缀， 限制此类下的所有方法请求url必须以请求前缀开头
3. 请求方法限定

- GET方法

``` @RequestMapping(method = RequestMethod.GET) ``` 

  - 如果通过POST访问则报错：
HTTP Status 405 - Request method 'POST' not supported

- POST方法

``` @RequestMapping(method = RequestMethod.POST) ``` 

  - 如果通过GET访问则报错：
HTTP Status 405 - Request method 'GET' not supported

## Controller 方法返回值

1. 返回ModelAndView
> 数据和视图
2. 返回String

```
@RequestMapping(value = "/item/itemlist.action")
    public String itemList(Model model) throws MessageException {

        // 从mysql中查询数据
        List<Items> list = itemService.selectItemsList();

        model.addAttribute("itemList",list);
        return "itemList";
    }
```

3. 返回void
> 在形参上可以定义request和response，使用request和response指定响应结果：

    1、使用request转发页面，如下：
    request.getRequestDispatcher("页面路径").forward(request, response);
    request.getRequestDispatcher("/WEB-INF/jsp/success.jsp").forward(request, response);
    
    2、可以通过response页面重定向：
    response.sendRedirect("url")
    response.sendRedirect("/springmvc-web2/itemEdit.action");
    
    3、可以通过response指定响应结果，例如响应json数据如下：
    response.getWriter().print("{\"abc\":123}");

## 异常处理
> 分为预期异常和运行时异常

![HandlerExceptionResolver](/img/HandlerExceptionResolver.png)

### 自定义异常处理器

实现HandlerExceptionResolver

```
/**
 * 异常处理器的自定义的实现类
 * Created by Jason on 2017/10/25.
 */
public class CustomExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object o, Exception e) {

        ModelAndView mav = new ModelAndView();
        //判断异常为类型
        if (e instanceof MessageException) {
            //预期异常
            mav.addObject("error",((MessageException) e).getMsg());

        } else {
            mav.addObject("error","未知异常");
        }
        mav.setViewName("error");
        return mav;

    }
}
```

## 上传图片

配置springmvc.xml

```
<!-- 文件上传,id必须设置为multipartResolver -->
<bean id="multipartResolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	<!-- 设置文件上传大小 -->
	<property name="maxUploadSize" value="5000000" />
</bean>
```

jsp页面，form表单设置

```
<form id="itemForm" action="${pageContext.request.contextPath }/updateitem.action" method="post" enctype="multipart/form-data">
    ......
</form>
```

controller添加上传图片

```
 //提交修改页面 入参为Items
    @RequestMapping(value = "/updateitem.action")
    public String updateitem(Items items, MultipartFile pictureFile) throws IOException {

        // 图片上传
        // 设置图片名称，不能重复，可以使用uuid
        String picName = UUID.randomUUID().toString();

        // 获取文件名
        String oriName = pictureFile.getOriginalFilename();
        // 获取图片后缀
        String extName = oriName.substring(oriName.lastIndexOf("."));

        // 开始上传
        pictureFile.transferTo(new File("D:/upload/" + picName + extName));


        items.setPic(picName + extName);

        itemService.updateItemsById(items);
        return "forward:/itemEdit.action";
    }
```

## json数据交互

### @RequestBody

作用：
@RequestBody注解用于读取**http请求的内容**(字符串)，通过springmvc提供的HttpMessageConverter接口将读到的内容（json数据）转换为java对象并绑定到Controller方法的参数上。

- 传统的请求参数：
itemEdit.action?id=1&name=zhangsan&age=12
- 现在的请求参数：
使用POST请求，在请求体里面加入json数据

```
{
"id": 1,
"name": "测试商品",
"price": 99.9,
"detail": "测试商品描述",
"pic": "123456.jpg"
}
```

### @ResponseBody

作用：
@ResponseBody注解用于将Controller的方法**返回的对象**，通过springmvc提供的HttpMessageConverter接口转换为指定格式的数据如：json,xml等，通过Response响应给客户端

```
    //json数据交互
    @RequestMapping(value = "/json.action")
    public @ResponseBody
    Items json(@RequestBody Items items) {
        return items;
    }
```

## 拦截器

> Spring Web MVC 的处理器拦截器类似于Servlet 开发中的过滤器Filter，用于对处理器进行预处理和后处理。

实现HandlerInterceptor

```
public class Interceptor1 implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object o) throws Exception {
        System.out.println("方法前1");
        //判断用户是否登录，如果没有，重定向到登录页面 不放行
        // URL http://localhost:8080/login.action
        // URI /login.action

        // 从request中获取session
        HttpSession session = request.getSession();
        // 从session中获取username
        Object username = session.getAttribute("username");
        // 判断username是否为null
        if (username != null) {
            // 如果不为空则放行
            return true;
        } else {
            // 如果为空则跳转到登录页面
            response.sendRedirect(request.getContextPath() + "/toLogin.action");
        }

        return false;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {
        System.out.println("方法后1");
    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        System.out.println("页面渲染后1");
    }
}
```

配置拦截器

```
<!-- 配置拦截器 -->
<mvc:interceptors>
	<mvc:interceptor>
		<!-- 所有的请求都进入拦截器 -->
		<mvc:mapping path="/**" />
		<!-- 配置具体的拦截器 -->
		<bean class="cn.itcast.ssm.interceptor.HandlerInterceptor1" />
	</mvc:interceptor>
</mvc:interceptors>
```

拦截器运行流程总结：
- preHandle按拦截器定义顺序调用
- postHandler按拦截器定义逆序调用
- afterCompletion按拦截器定义逆序调用
- postHandler在拦截器链内所有拦截器返成功调用
- afterCompletion只有preHandle返回true才调用


