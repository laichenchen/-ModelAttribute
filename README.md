# -ModelAttribute
具体使用及作用

1.@ModelAttribute注释void返回值的方法
@Controller  
public class HelloModelController {  
      
    @ModelAttribute   
    public void populateModel(@RequestParam String abc, Model model) {    
       model.addAttribute("attributeName", abc);    
    }    
  
    @RequestMapping(value = "/helloWorld")    
    public String helloWorld() {    
       return "helloWorld.jsp";    
    }    
  
}  
在这个代码中，访问控制器方法helloWorld时，会首先调用populateModel方法，
将页面参数abc(/helloWorld.html?abc=text)放到model的attributeName属性中，在视图中可以直接访问。

jsp页面页面如下：
<c:out value="${attributeName}"></c:out>  

2.@ModelAttribute注释返回具体类的方法
@Controller  
public class Hello2ModelController {  
      
    @ModelAttribute   
    public User populateModel() {    
       User user=new User();  
       user.setAccount("ray");  
       return user;  
    }    
    @RequestMapping(value = "/helloWorld2")    
    public String helloWorld() {    
       return "helloWorld.jsp";    
    }    
}  
当用户请求 http://localhost:8080/test/helloWorld2.html时，
首先访问populateModel方法，返回User对象，model属性的名称没有指定，它由返回类型隐含表示，
如这个方法返回User类型，那么这个model属性的名称是user。 
这个例子中model属性名称有返回对象类型隐含表示，model属性对象就是方法的返回值。它无须要特定的参数。

jsp 中如下访问：
<c:out value="${user.account}"></c:out>  

也可以指定属性名称
@Controller  
public class Hello2ModelController {  
      
    @ModelAttribute(value="myUser")  
    public User populateModel() {    
       User user=new User();  
       user.setAccount("ray");  
       return user;  
    }    
    @RequestMapping(value = "/helloWorld2")    
    public String helloWorld(Model map) {    
       return "helloWorld.jsp";    
    }    
}  
jsp中如下访问：
<c:out value="${myUser.account}"></c:out>  


总结：
一、绑定请求参数到指定对象     
 
public String test1(@ModelAttribute("user") UserModel user)  
 参数中多了一个注解@ModelAttribute("user").
 它的作用是将该绑定的命令对象以“user”为名称添加到模型对象中供视图页面展示使用。
 我们此时可以在视图页面使用${user.username}来获取绑定的命令对象的属性。
 
二、暴露表单引用对象为模型数据 
通过这个方法可以显示出一个集合的内容
/**
 * 设置这个注解之后可以直接在前端页面使用hb这个对象（List）集合
 * @return
 */
@ModelAttribute("hb")
public List<String> hobbiesList(){
	List<String> hobbise = new LinkedList<String>();
	hobbise.add("basketball");
	hobbise.add("football");
	hobbise.add("tennis");
	return hobbise;
}

<br>
初始化的数据 ： 	${hb }
<br>

	<c:forEach items="${hb}" var="hobby" varStatus="vs">
		<c:choose>
			<c:when test="${hobby == 'basketball'}">
			篮球<input type="checkbox" name="hobbies" value="basketball">
			</c:when>
			<c:when test="${hobby == 'football'}">
				足球<input type="checkbox" name="hobbies" value="football">
			</c:when>
			<c:when test="${hobby == 'tennis'}">
				网球<input type="checkbox" name="hobbies" value="tennis">
			</c:when>
		</c:choose>
	</c:forEach>
  
 三、暴露@RequestMapping方法返回值为模型数据 
 public @ModelAttribute("user2") UserModel test3(@ModelAttribute("user2") UserModel user)
 
  
  
