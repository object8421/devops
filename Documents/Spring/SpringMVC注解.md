# Spring MVC注解详解 [4.x]

## @Controller

该注解使用在控制层的类上，表明该类是一个controller容器。

### 参数

* `name`: 可选。设置控制器的名称，通常为系统默认（类名的首字母小字）

## @RequestMapping

请求路径映射，如果标注在某个controller的类级别上，则表明访问此类路径下的方法都要加上其配置的路径；最常用是标注在方法上，表明哪个具体的方法来接受处理某次请求。

### 参数

* `value`：指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）。

* `method`：  指定请求的method类型， GET、POST、PUT、DELETE等；

* `consumes`： 指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;

* `produces`: 指定返回的内容类型，仅当request请求头的(Accept)类型中包含该指定类型才返回；

* `params`： 指定request中必须包含某些参数值是，才让该方法处理。

* `headers`： 指定request中必须包含某些指定的header值，才能让该方法处理请求。

## @PathVariable 

当使用@RequestMapping URI template 样式映射时， 即 someUrl/{paramId}, 这时的paramId可通过 @Pathvariable注解绑定它传过来的值到方法的参数上。

```
@Controller
@RequestMapping("/owners/{ownerId}")
public class RelativePathUriTemplateController {

  @RequestMapping("/pets/{petId}")
  public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {    
    // implementation omitted
  }
}
```

_Spring 3.x中使用@PathVariable时必须指定名称值，如果名称相同可以不指定，该BUG在Spring 4.x中已修复_

## @RequestHeader

@RequestHeader 注解，可以把Request请求header部分的值绑定到方法的参数上。@RequestHeader 注解，可以把Request请求header部分的值绑定到方法的参数上。

```
这是一个Request 的header部分：
Host                    localhost:8080
Accept                  text/html,application/xhtml+xml,application/xml;q=0.9
Accept-Language         fr,en-gb;q=0.7,en;q=0.3
Accept-Encoding         gzip,deflate
Accept-Charset          ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive              300

@RequestMapping("/displayHeaderInfo.do")
public void displayHeaderInfo(@RequestHeader("Accept-Encoding") String encoding, @RequestHeader("Keep-Alive") long keepAlive)  {
  //...
}
```

## @CookieValue

@CookieValue 可以把Request header中关于cookie的值绑定到方法的参数上。

```
例如有如下Cookie值： JSESSIONID=415A4AC178C59DACE0B2C9CA727CDD84

参数绑定的代码：
@RequestMapping("/displayHeaderInfo.do")
public void displayHeaderInfo(@CookieValue("JSESSIONID") String cookie)  {
  //...
}
即把JSESSIONID的值绑定到参数cookie上。
```

## @RequestParam

* A） 常用来处理简单类型的绑定，通过Request.getParameter() 获取的String可直接转换为简单类型的情况（ String--> 简单类型的转换操作由ConversionService配置的转换器来完成）；因为使用request.getParameter()方式获取参数，所以可以处理get 方式中queryString的值，也可以处理post方式中 body data的值；

* B）用来处理Content-Type: 为 application/x-www-form-urlencoded编码的内容，提交方式GET、POST；

* C) 该注解有两个属性： value、required； value用来指定要传入值的id名称，required用来指示参数是否必须绑定；

```
@Controller
@RequestMapping("/pets")
@SessionAttributes("pet")
public class EditPetForm {
    @RequestMapping(method = RequestMethod.GET)
    public String setupForm(@RequestParam("petId") int petId, ModelMap model) {
        Pet pet = this.clinic.loadPet(petId);
        model.addAttribute("pet", pet);
        return "petForm";
    }
```

## @RequestBody

该注解常用来处理Content-Type: 不是application/x-www-form-urlencoded编码的内容，例如application/json, application/xml等；

它是通过使用HandlerAdapter 配置的HttpMessageConverters来解析post data body，然后绑定到相应的bean上的。

因为配置有FormHttpMessageConverter，所以也可以用来处理 application/x-www-form-urlencoded的内容，处理完的结果放在一个MultiValueMap<String, String>里，这种情况在某些特殊需求下使用，详情查看FormHttpMessageConverter api;

```
@RequestMapping(value = "/something", method = RequestMethod.PUT)
public void handle(@RequestBody String body, Writer writer) throws IOException {
  writer.write(body);
}
```

## @SessionAttributes

该注解用来绑定HttpSession中的attribute对象的值，便于在方法中的参数里使用。

该注解有value、types两个属性，可以通过名字和类型指定要使用的attribute 对象；

```
@Controller
@RequestMapping("/editPet.do")
@SessionAttributes("pet")
public class EditPetForm {
    // ...
}
```

## @ModelAttribute

该注解有两个用法，一个是用于方法上，一个是用于参数上；

用于方法上时：  通常用来在处理@RequestMapping之前，为请求绑定需要从后台查询的model；

用于参数上时： 用来通过名称对应，把相应名称的值绑定到注解的参数bean上；要绑定的值来源于：

A） @SessionAttributes 启用的attribute 对象上；

B） @ModelAttribute 用于方法上时指定的model对象；

C） 上述两种情况都没有时，new一个需要绑定的bean对象，然后把request中按名称对应的方式把值绑定到bean中。

```
用到方法上@ModelAttribute的示例代码：
// Add one attribute
// The return value of the method is added to the model under the name "account"
// You can customize the name via @ModelAttribute("myAccount")

@ModelAttribute
public Account addAccount(@RequestParam String number) {
    return accountManager.findAccount(number);
}
这种方式实际的效果就是在调用@RequestMapping的方法之前，为request对象的model里put（“account”， Account）；

用在参数上的@ModelAttribute示例代码：

@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)
public String processSubmit(@ModelAttribute Pet pet) {
   
}
首先查询 @SessionAttributes有无绑定的Pet对象，若没有则查询@ModelAttribute方法层面上是否绑定了Pet对象，若没有则将URI template中的值按对应的名称绑定到Pet对象的各属性上。
```