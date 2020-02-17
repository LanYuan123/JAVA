# SpringMVC常用注解

## :palm_tree:@RequestMapping

   RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上，用在类上时，该注解配置的URI是所有该类方法的URI的前缀
   
   @Request有六个属性：
   
   ### value
   
   指定请求的URI地址
   
   URI的值可以为以下三类
   - 普通的具体值
   ```
    @RequestMapping(value="/new")
    public AppointmentForm getNewForm() {
        return new AppointmentForm();
    }

   ```
   - 含有某变量的一类值，和@PathVariable注解一起使用
   ```
    @RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
    public String findOwner(@PathVariable String ownerId, Model model) {
      Owner owner = ownerService.findOwner(ownerId);  
      model.addAttribute("owner", owner);  
      return "displayOwner"; 
    }
   ```
   - 可以指定为含正则表达式的一类值，和@PathVariable注解一起使用
   ```
   @RequestMapping("/spring-web/{symbolicName:[a-z-]+}-{version:\d\.\d\.\d}.{extension:\.[a-z]}")
     public void handle(@PathVariable String version, @PathVariable String extension) {    
       // ...
     }
   }
   ```
   
   ### method
   
   指定请求的method类型，GET、POST、PUT、DELETE等
   ```
    @RequestMapping(method = RequestMethod.GET)
    public Map<String, Appointment> get() {
        return appointmentBook.getAppointmentsForToday();
    }
   ```
   
   ### consumes
   
   指定处理请求的提交内容类型（Content-Type），例如application/json,text/html
   ```
    @Controller
    @RequestMapping(value = "/pets", method = RequestMethod.POST, consumes="application/json")
    public void addPet(@RequestBody Pet pet, Model model) {    
        // implementation omitted
    }
    
    方法仅处理request Content-Type为“application/json”类型的请求
   ```
   
   ### produces
   
   指定返回的内容类型，仅当request请求头中的（Accept）类型中包含该指定类型才返回
   ```
    @Controller
    @RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, produces="application/json")
    @ResponseBody
    public Pet getPet(@PathVariable String petId, Model model) {    
        // implementation omitted
    }
    
    方法仅处理request请求中Accept头中包含了"application/json"的请求，同时暗示了返回的内容类型为application/json;
   ```
   
   ### params
   
   指定request中必须包含某些参数值，才让该方法处理
   ```
    @Controller
    @RequestMapping("/owners/{ownerId}")
    public class RelativePathUriTemplateController {

      @RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, params="myParam=myValue")
      public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {    
        // implementation omitted
      }
    }
    
    仅处理请求中包含了名为“myParam”，值为“myValue”的请求；
   ```
   
   ### headers
   
   指定request中必须包含某些指定的header值，才能让该方法处理请求
   ```
    @Controller
    @RequestMapping("/owners/{ownerId}")
    public class RelativePathUriTemplateController {

    @RequestMapping(value = "/pets", method = RequestMethod.GET, headers="Referer=http://www.ifeng.com/")
      public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {    
        // implementation omitted
      }
    }
    
    仅处理request的header中包含了指定“Refer”请求头和对应值为“http://www.ifeng.com/”的请求；
   ```

## :palm_tree:@RequestParam

   当方法中的参数和请求参数类型，名称一致的时候，会自动匹配参数到方法所需的参数之中，但是当方法中的参数名称和请求参数的名称不匹配的时候，那么使用@RequestParam注解去绑定请求参数值，@RequestParam只能处理Content-Type类型为application/x-www-form-urlencoded的参数
   
   @RequestParam注解有三个属性：
   ### value
   
   请求参数的参数名
   
   ### required
   
   该参数是否必须，默认为true
   
   ### defaultValue
   
   请求参数的默认值，表示如果请求参数中没有该参数，则使用defaultValue设置的默认值，设置值的时候必须是String，框架会自动转换

## :palm_tree:@RequestBody

   该注解用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上，再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上。 

## @RequestParam和@RequestBody之间的区别


## :palm_tree:@ResponseBody

## :palm_tree:@RestController

## :palm_tree:@PathVariable

## :palm_tree:@RequestHeader

## :palm_tree:@CookieValue

## :palm_tree:@GetMapping

## :palm_tree:@PostMapping

## :palm_tree:@DeleteMapping

## :palm_tree:@ModelAttribute

## :palm_tree:@SessionAttribute


> 参考文章：[@RequestMapping 用法详解之地址映射](https://blog.csdn.net/walkerJong/article/details/7994326)
