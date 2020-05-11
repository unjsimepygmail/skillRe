# spring review

## Factorybean和beanfactory有什么区别
> + beanfactory是spring的IOC的容器，用于管理bean的一个工厂
> + beanfactory是一个接口，常用实现类：ApplicationContext/xmlBeanfactory/DefaultListableBeanFactory
> + factorybean是一个特殊的bean，能生产或者修饰对象生成的工厂bean，它的实现与设计模式中的工厂模式和修饰器模式类似。

## 引入装饰器模式
> + 意图：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
> + 主要解决：一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。

## spring IOC
> + IOC控制反转是一种思想，将原本需要手动管理的对象统一由spring的IOC容器去管理，使用时可以通过注解的方式去获取对象，不需要在任何service中去new对象。
* IOC注入几种方式

> + 接口注入：
接口注入模式因为具备侵入性，它要求组件必须与特定的接口相关联，因此并不被看好，实际使用有限。


> + Setter 注入：
对于习惯了传统 javabean 开发的程序员，通过 setter 方法设定依赖关系更加直观。如果依赖关系较为复杂，那么构造子注入模式的构造函数也会相当庞大，而此时设值注入模式则更为简洁。如果用到了第三方类库，可能要求我们的组件提供一个默认的构造函数，此时构造子注入模式也不适用。


> + 构造器注入：
在构造期间完成一个完整的、合法的对象。所有依赖关系在构造函数中集中呈现。依赖关系在构造时由容器一次性设定，组件被创建之后一直处于相对“不变”的稳定状态。只有组件的创建者关心其内部依赖关系，对调用者而言，该依赖关系处于“黑盒”之中。

## spring bean的生命周期

> + beanfactory通过反射??去实例化一个bean
> + 填充bean的属性 pring上下文对实例化的Bean进行配置－－也就是IOC注入
> + 如果bean实现了BeanNameAware接口，则会调用他的SetBeanName(Str)函数，此处的str就是定义的bean的id
> + 如果bean实现了BeanFactoryAware接口，会调用它的SetBeanFactory方法，参数是spring工厂自身
> + 如果实现了ApplicationContextAware接口 则会调用setApplicationContext(ApplicationContext)传入spring上下文，同样这个方式也可以实现步骤4的内容，但比4更好，因为ApplicationContext是BeanFactory的子接口，有更多的实现方法）；
> + 如果Bean实现了BeanPostProcessor接口，则调用其postProcessBeforeInitialization方法。
> + 如果Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法
> + 如果有和Bean关联的BeanPostProcessors对象，则这些对象的postProcessAfterInitialization方法被调用。
> + 当销毁Bean实例时，如果Bean实现了DisposableBean接口，则调用其destroy方法。
> + 最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。

## Spring bean的作用域

> + singleton：默认的scope，每个scope为singleton的bean都会被定义为一个单例对象，该对象的生命周期是与Spring IOC容器一致的（但在第一次被注入时才会创建）。

> + prototype：bean被定义为在每次注入时都会创建一个新的对象。

> + request：bean被定义为在每个HTTP请求中创建一个单例对象，也就是说在单个请求中都会复用这一个单例对象。

> + session：bean被定义为在一个session的生命周期内创建一个单例对象。

> + application：bean被定义为在ServletContext的生命周期中复用一个单例对象。

> + websocket：bean被定义为在websocket的生命周期中复用一个单例对象。

## 引入浅拷贝和深拷贝

> + 浅拷贝：比如一个对象中含有引用类型，则引用类型不会被拷贝一份。
> + 深拷贝: 对象中的所有类型都会被在堆内存中拷贝一份
